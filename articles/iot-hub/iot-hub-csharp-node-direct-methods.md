---
title: aaaUse Azure IoT Hub direkt metoder (.NET/nod) | Microsoft Docs
description: "Hur toouse Azure IoT Hub direkt metoder. Du kan använda hello Azure IoT-enhet SDK för Node.js tooimplement en simulerad enhetsapp som innehåller en direkt metod och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som anropar hello direkta metoden."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: nberdy
ms.openlocfilehash: f566f939be840eb308b00ffa4e05c4e5b3fefb39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-netnode"></a><span data-ttu-id="e42b8-104">Använda direkt metoder (.NET/nod)</span><span class="sxs-lookup"><span data-stu-id="e42b8-104">Use direct methods (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="e42b8-105">I den här självstudiekursen kommer vi toodevelop en .NET och Node.js-konsolapp:</span><span class="sxs-lookup"><span data-stu-id="e42b8-105">In this tutorial, we are going toodevelop a .NET and a Node.js console app:</span></span>

* <span data-ttu-id="e42b8-106">**CallMethodOnDevice.sln**, en .NET backend-app, som anropar en metod i hello simulerade enhetsapp och visar hello svar.</span><span class="sxs-lookup"><span data-stu-id="e42b8-106">**CallMethodOnDevice.sln**, a .NET back-end app, which calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="e42b8-107">**SimulatedDevice.js**, en Node.js-app som simulerar en enhet som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och svarar toohello-metoden anropas av hello molnet.</span><span class="sxs-lookup"><span data-stu-id="e42b8-107">**SimulatedDevice.js**, a Node.js app, which simulates a device connecting tooyour IoT hub with hello device identity created earlier, and responds toohello method called by hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="e42b8-108">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild toorun både program på enheter och din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="e42b8-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="e42b8-109">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="e42b8-109">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="e42b8-110">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e42b8-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="e42b8-111">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="e42b8-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="e42b8-112">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e42b8-112">An active Azure account.</span></span> <span data-ttu-id="e42b8-113">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="e42b8-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="e42b8-114">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="e42b8-114">Create a simulated device app</span></span>
<span data-ttu-id="e42b8-115">I det här avsnittet skapar du en Node.js-konsolprogram som svarar tooa metoden anropas av hello-lösningen slutet.</span><span class="sxs-lookup"><span data-stu-id="e42b8-115">In this section, you create a Node.js console app that responds tooa method called by hello solution back end.</span></span>

1. <span data-ttu-id="e42b8-116">Skapa en ny tom mapp med namnet **simulateddevice**.</span><span class="sxs-lookup"><span data-stu-id="e42b8-116">Create a new empty folder called **simulateddevice**.</span></span> <span data-ttu-id="e42b8-117">I hello **simulateddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="e42b8-117">In hello **simulateddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="e42b8-118">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="e42b8-118">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="e42b8-119">Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** och **azure-iot-enhet – mqtt** paket:</span><span class="sxs-lookup"><span data-stu-id="e42b8-119">At your command prompt in hello **simulateddevice** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="e42b8-120">Med hjälp av en textredigerare, skapa en fil i hello **simulateddevice** mapp och ger den namnet **SimulatedDevice.js**.</span><span class="sxs-lookup"><span data-stu-id="e42b8-120">Using a text editor, create a file in hello **simulateddevice** folder and name it **SimulatedDevice.js**.</span></span>
4. <span data-ttu-id="e42b8-121">Lägg till följande hello `require` uttryck högst hello början av hello **SimulatedDevice.js** fil:</span><span class="sxs-lookup"><span data-stu-id="e42b8-121">Add hello following `require` statements at hello start of hello **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. <span data-ttu-id="e42b8-122">Lägg till en **connectionString** variabel och använda den toocreate en **DeviceClient** instans.</span><span class="sxs-lookup"><span data-stu-id="e42b8-122">Add a **connectionString** variable and use it toocreate a **DeviceClient** instance.</span></span> <span data-ttu-id="e42b8-123">Ersätt **{enheten anslutningssträngen}** med hello enheten anslutningssträng som du genererade i hello *skapa en enhetsidentitet* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="e42b8-123">Replace **{device connection string}** with hello device connection string you generated in hello *Create a device identity* section:</span></span>
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. <span data-ttu-id="e42b8-124">Lägg till hello följande funktion tooimplement hello direkt metod på hello enhet:</span><span class="sxs-lookup"><span data-stu-id="e42b8-124">Add hello following function tooimplement hello direct method on hello device:</span></span>
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written toolog.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. <span data-ttu-id="e42b8-125">Öppna hello anslutning tooyour IoT-hubb och initiera lyssnaren för hello-metoden:</span><span class="sxs-lookup"><span data-stu-id="e42b8-125">Open hello connection tooyour IoT hub and initialize hello method listener:</span></span>
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. <span data-ttu-id="e42b8-126">Spara och Stäng hello **SimulatedDevice.js** fil.</span><span class="sxs-lookup"><span data-stu-id="e42b8-126">Save and close hello **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="e42b8-127">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="e42b8-127">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="e42b8-128">I produktionskod, bör du implementera försök principer (till exempel anslutning försök), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="e42b8-128">In production code, you should implement retry policies (such as connection retry), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="e42b8-129">Anropa en metod som direkt på en enhet</span><span class="sxs-lookup"><span data-stu-id="e42b8-129">Call a direct method on a device</span></span>
<span data-ttu-id="e42b8-130">I det här avsnittet skapar du en .NET-konsolapp som anropar en metod i hello simulerade enheten appen och sedan visar hello svar.</span><span class="sxs-lookup"><span data-stu-id="e42b8-130">In this section, you create a .NET console app that calls a method in hello simulated device app and then displays hello response.</span></span>

1. <span data-ttu-id="e42b8-131">I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="e42b8-131">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="e42b8-132">Se till att hello av .NET Framework 4.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e42b8-132">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="e42b8-133">Namnet hello projektet **CallMethodOnDevice**.</span><span class="sxs-lookup"><span data-stu-id="e42b8-133">Name hello project **CallMethodOnDevice**.</span></span>
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][10]
2. <span data-ttu-id="e42b8-135">I Solution Explorer högerklickar du på hello **CallMethodOnDevice** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="e42b8-135">In Solution Explorer, right-click hello **CallMethodOnDevice** project, and then click **Manage NuGet Packages...**.</span></span>
3. <span data-ttu-id="e42b8-136">I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="e42b8-136">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="e42b8-137">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="e42b8-137">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Fönstret för NuGet-pakethanteraren][11]

4. <span data-ttu-id="e42b8-139">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="e42b8-139">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="e42b8-140">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="e42b8-140">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="e42b8-141">Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="e42b8-141">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="e42b8-142">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="e42b8-142">Add hello following method toohello **Program** class:</span></span>
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line toobe written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    <span data-ttu-id="e42b8-143">Den här metoden startar direkt metod med namnet `writeLine` på hello `myDeviceId` enhet.</span><span class="sxs-lookup"><span data-stu-id="e42b8-143">This method invokes a direct method with name `writeLine` on hello `myDeviceId` device.</span></span> <span data-ttu-id="e42b8-144">Sedan skriver hello-svar som tillhandahålls av hello enheten på hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="e42b8-144">Then, it writes hello response provided by hello device on hello console.</span></span> <span data-ttu-id="e42b8-145">Observera hur det är möjligt toospecify ett timeout-värde för hello enheten toorespond.</span><span class="sxs-lookup"><span data-stu-id="e42b8-145">Note how it is possible toospecify a timeout value for hello device toorespond.</span></span>
7. <span data-ttu-id="e42b8-146">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="e42b8-146">Finally, add hello following lines toohello **Main** method:</span></span>
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

## <a name="run-hello-applications"></a><span data-ttu-id="e42b8-147">Köra hello program</span><span class="sxs-lookup"><span data-stu-id="e42b8-147">Run hello applications</span></span>
<span data-ttu-id="e42b8-148">Du är nu redo toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="e42b8-148">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="e42b8-149">I hello Visual Studio Solution Explorer högerklickar du på din lösning och klicka sedan på **ange Startprojekt...** . Välj **enda Startprojekt**, och välj sedan hello **CallMethodOnDevice** projekt i hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="e42b8-149">In hello Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**. Select **Single startup project**, and then select hello **CallMethodOnDevice** project in hello dropdown menu.</span></span>

2. <span data-ttu-id="e42b8-150">Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando toostart lyssnar efter metodanrop från din IoT-hubb hello:</span><span class="sxs-lookup"><span data-stu-id="e42b8-150">At a command prompt in hello **simulateddevice** folder, run hello following command toostart listening for method calls from your IoT Hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   <span data-ttu-id="e42b8-151">Vänta tills hello simulerade enheten tooopen:![][7]</span><span class="sxs-lookup"><span data-stu-id="e42b8-151">Wait for hello simulated device tooopen:  ![][7]</span></span>
3. <span data-ttu-id="e42b8-152">Nu hello enheten är ansluten och väntar på att metoden anrop, kör hello .NET **CallMethodOnDevice** tooinvoke hello mobilappmetoden i hello simulerade enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="e42b8-152">Now that hello device is connected and waiting for method invocations, run hello .NET **CallMethodOnDevice** app tooinvoke hello method in hello simulated device app.</span></span> <span data-ttu-id="e42b8-153">Du bör se hello enhetens svar skrivs i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="e42b8-153">You should see hello device response written in hello console.</span></span>
   
    ![][8]
4. <span data-ttu-id="e42b8-154">hello enheten reagerar sedan toohello metoden genom att skriva ut det här meddelandet:</span><span class="sxs-lookup"><span data-stu-id="e42b8-154">hello device then reacts toohello method by printing this message:</span></span>
   
    ![][9]

## <a name="next-steps"></a><span data-ttu-id="e42b8-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e42b8-155">Next steps</span></span>
<span data-ttu-id="e42b8-156">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="e42b8-156">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="e42b8-157">Du har använt den här enheten identitet tooenable hello simulerade enheten app tooreact toomethods anropas av hello molnet.</span><span class="sxs-lookup"><span data-stu-id="e42b8-157">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="e42b8-158">Skapade du även en app som anropar metoder på hello enhet och visar hello svar från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="e42b8-158">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span> 

<span data-ttu-id="e42b8-159">toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:</span><span class="sxs-lookup"><span data-stu-id="e42b8-159">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="e42b8-160">[Kom igång med IoT-hubb]</span><span class="sxs-lookup"><span data-stu-id="e42b8-160">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="e42b8-161">[Schema-jobb på flera enheter][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="e42b8-161">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="e42b8-162">toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="e42b8-162">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

<!-- Images. -->
[7]: ./media/iot-hub-csharp-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-csharp-node-direct-methods/netserviceapp.png
[9]: ./media/iot-hub-csharp-node-direct-methods/methods-output.png

[10]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp1.png
[11]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp2.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Kom igång med IoT-hubb]: iot-hub-node-node-getstarted.md
