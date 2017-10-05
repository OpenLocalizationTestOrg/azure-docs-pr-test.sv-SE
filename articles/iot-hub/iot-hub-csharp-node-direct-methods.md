---
title: "Använda Azure IoT Hub direkt metoder (.NET/nod) | Microsoft Docs"
description: "Hur du använder Azure IoT Hub direkt metoder. Du kan använda Azure IoT-enhet SDK för Node.js för att implementera en simulerad enhetsapp som innehåller en direkt metod och Azure IoT SDK för .NET att implementera ett service-appen som anropar metoden direkt."
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
ms.openlocfilehash: ad705789a153381e21b2ccb05d4e0c17f78671fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-direct-methods-netnode"></a><span data-ttu-id="11b3e-104">Använda direkt metoder (.NET/nod)</span><span class="sxs-lookup"><span data-stu-id="11b3e-104">Use direct methods (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="11b3e-105">I den här kursen ska vi ta fram en .NET och Node.js-konsolapp:</span><span class="sxs-lookup"><span data-stu-id="11b3e-105">In this tutorial, we are going to develop a .NET and a Node.js console app:</span></span>

* <span data-ttu-id="11b3e-106">**CallMethodOnDevice.sln**, en .NET backend-app, som anropar en metod i appen simulerade enheten och visar svaret.</span><span class="sxs-lookup"><span data-stu-id="11b3e-106">**CallMethodOnDevice.sln**, a .NET back-end app, which calls a method in the simulated device app and displays the response.</span></span>
* <span data-ttu-id="11b3e-107">**SimulatedDevice.js**, en Node.js-app som simulerar en enhet som ansluter till din IoT-hubb med enhetens identitet skapade tidigare och svarar på metoden som anropades av molnet.</span><span class="sxs-lookup"><span data-stu-id="11b3e-107">**SimulatedDevice.js**, a Node.js app, which simulates a device connecting to your IoT hub with the device identity created earlier, and responds to the method called by the cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="11b3e-108">Artikeln om [Azure IoT SDK:er][lnk-hub-sdks] innehåller information om Azure IoT SDK:er som du kan använda för att skapa båda apparna så att de kan köras på enheter och på lösningens backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="11b3e-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both applications to run on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="11b3e-109">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="11b3e-109">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="11b3e-110">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="11b3e-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="11b3e-111">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="11b3e-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="11b3e-112">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="11b3e-112">An active Azure account.</span></span> <span data-ttu-id="11b3e-113">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="11b3e-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="11b3e-114">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="11b3e-114">Create a simulated device app</span></span>
<span data-ttu-id="11b3e-115">I det här avsnittet skapar du en Node.js-konsolprogram som svarar på en metod som anropas av lösningen tillbaka slutet.</span><span class="sxs-lookup"><span data-stu-id="11b3e-115">In this section, you create a Node.js console app that responds to a method called by the solution back end.</span></span>

1. <span data-ttu-id="11b3e-116">Skapa en ny tom mapp med namnet **simulateddevice**.</span><span class="sxs-lookup"><span data-stu-id="11b3e-116">Create a new empty folder called **simulateddevice**.</span></span> <span data-ttu-id="11b3e-117">I mappen **simulateddevice** skapar du en package.json-fil med hjälp av följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="11b3e-117">In the **simulateddevice** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="11b3e-118">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="11b3e-118">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="11b3e-119">Vid en kommandotolk i den **simulateddevice** mapp, kör följande kommando för att installera den **azure iot-enhet** och **azure-iot-enhet – mqtt** paket:</span><span class="sxs-lookup"><span data-stu-id="11b3e-119">At your command prompt in the **simulateddevice** folder, run the following command to install the **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="11b3e-120">Med hjälp av en textredigerare, skapa en fil i den **simulateddevice** mapp och ger den namnet **SimulatedDevice.js**.</span><span class="sxs-lookup"><span data-stu-id="11b3e-120">Using a text editor, create a file in the **simulateddevice** folder and name it **SimulatedDevice.js**.</span></span>
4. <span data-ttu-id="11b3e-121">Lägg till följande `require`-instruktioner i början av **SimulatedDevice.js**-filen:</span><span class="sxs-lookup"><span data-stu-id="11b3e-121">Add the following `require` statements at the start of the **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. <span data-ttu-id="11b3e-122">Lägg till en **connectionString** variabel och använda den för att skapa en **DeviceClient** instans.</span><span class="sxs-lookup"><span data-stu-id="11b3e-122">Add a **connectionString** variable and use it to create a **DeviceClient** instance.</span></span> <span data-ttu-id="11b3e-123">Ersätt **{enheten anslutningssträngen}** med enhetsanslutningen sträng som du genererade i den *skapa en enhetsidentitet* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="11b3e-123">Replace **{device connection string}** with the device connection string you generated in the *Create a device identity* section:</span></span>
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. <span data-ttu-id="11b3e-124">Lägg till följande funktion för att implementera metoden direkt på enheten:</span><span class="sxs-lookup"><span data-stu-id="11b3e-124">Add the following function to implement the direct method on the device:</span></span>
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. <span data-ttu-id="11b3e-125">Öppna anslutning till din IoT-hubb och initiera lyssnaren för metoden:</span><span class="sxs-lookup"><span data-stu-id="11b3e-125">Open the connection to your IoT hub and initialize the method listener:</span></span>
   
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
8. <span data-ttu-id="11b3e-126">Spara och stäng filen **SimulatedDevice.js**.</span><span class="sxs-lookup"><span data-stu-id="11b3e-126">Save and close the **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="11b3e-127">För att göra det så enkelt som möjligt implementerar vi ingen princip för omförsök i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="11b3e-127">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="11b3e-128">I produktionskod, bör du implementera försök principer (till exempel anslutning försök), enligt förslaget i MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="11b3e-128">In production code, you should implement retry policies (such as connection retry), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="11b3e-129">Anropa en metod som direkt på en enhet</span><span class="sxs-lookup"><span data-stu-id="11b3e-129">Call a direct method on a device</span></span>
<span data-ttu-id="11b3e-130">I det här avsnittet skapar du en .NET-konsolapp som anropar en metod i appen simulerade enheten och sedan visar svaret.</span><span class="sxs-lookup"><span data-stu-id="11b3e-130">In this section, you create a .NET console app that calls a method in the simulated device app and then displays the response.</span></span>

1. <span data-ttu-id="11b3e-131">I Visual Studio lägger du till ett Visual C# Classic Desktop-projekt i den aktuella lösningen med hjälp av projektmallen **Konsolprogram**.</span><span class="sxs-lookup"><span data-stu-id="11b3e-131">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="11b3e-132">Kontrollera att .NET Framework-versionen är 4.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="11b3e-132">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="11b3e-133">Namnge projektet **CallMethodOnDevice**.</span><span class="sxs-lookup"><span data-stu-id="11b3e-133">Name the project **CallMethodOnDevice**.</span></span>
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][10]
2. <span data-ttu-id="11b3e-135">I Solution Explorer högerklickar du på den **CallMethodOnDevice** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="11b3e-135">In Solution Explorer, right-click the **CallMethodOnDevice** project, and then click **Manage NuGet Packages...**.</span></span>
3. <span data-ttu-id="11b3e-136">Välj **Bläddra** i fönstret för **NuGet-pakethanteraren**, leta upp **microsoft.azure.devices**, välj **Installera** för att installera **Microsoft.Azure.Devices**-paketet och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="11b3e-136">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="11b3e-137">Denna procedur hämtar, installerar och lägger till en referens för [NuGet-paketetet SDK för Azure IoT-tjänster][lnk-nuget-service-sdk] och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="11b3e-137">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Fönstret för NuGet-pakethanteraren][11]

4. <span data-ttu-id="11b3e-139">Lägg till följande `using`-uttryck överst i **Program.cs**-filen:</span><span class="sxs-lookup"><span data-stu-id="11b3e-139">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="11b3e-140">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="11b3e-140">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="11b3e-141">Ersätt platshållarvärdet med IoT Hub-anslutningssträngen som du skapade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="11b3e-141">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="11b3e-142">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="11b3e-142">Add the following method to the **Program** class:</span></span>
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line to be written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    <span data-ttu-id="11b3e-143">Den här metoden startar direkt metod med namnet `writeLine` på den `myDeviceId` enhet.</span><span class="sxs-lookup"><span data-stu-id="11b3e-143">This method invokes a direct method with name `writeLine` on the `myDeviceId` device.</span></span> <span data-ttu-id="11b3e-144">Sedan skriver svaret från enheten på konsolen.</span><span class="sxs-lookup"><span data-stu-id="11b3e-144">Then, it writes the response provided by the device on the console.</span></span> <span data-ttu-id="11b3e-145">Observera hur är det möjligt att ange en tidsgräns för enheten att svara.</span><span class="sxs-lookup"><span data-stu-id="11b3e-145">Note how it is possible to specify a timeout value for the device to respond.</span></span>
7. <span data-ttu-id="11b3e-146">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="11b3e-146">Finally, add the following lines to the **Main** method:</span></span>
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

## <a name="run-the-applications"></a><span data-ttu-id="11b3e-147">Köra programmen</span><span class="sxs-lookup"><span data-stu-id="11b3e-147">Run the applications</span></span>
<span data-ttu-id="11b3e-148">Nu är det dags att köra programmen.</span><span class="sxs-lookup"><span data-stu-id="11b3e-148">You are now ready to run the applications.</span></span>

1. <span data-ttu-id="11b3e-149">Högerklicka på lösningen i Visual Studio Solution Explorer och klicka sedan på **ange Startprojekt...** .</span><span class="sxs-lookup"><span data-stu-id="11b3e-149">In the Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**.</span></span> <span data-ttu-id="11b3e-150">Välj **enda Startprojekt**, och välj sedan den **CallMethodOnDevice** projekt i den nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="11b3e-150">Select **Single startup project**, and then select the **CallMethodOnDevice** project in the dropdown menu.</span></span>

2. <span data-ttu-id="11b3e-151">Vid en kommandotolk i den **simulateddevice** mapp, kör följande kommando för att påbörja avlyssning för metodanrop från din IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="11b3e-151">At a command prompt in the **simulateddevice** folder, run the following command to start listening for method calls from your IoT Hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   <span data-ttu-id="11b3e-152">Vänta tills den simulerade enheten att öppna:![][7]</span><span class="sxs-lookup"><span data-stu-id="11b3e-152">Wait for the simulated device to open:  ![][7]</span></span>
3. <span data-ttu-id="11b3e-153">Nu när enheten är ansluten och väntar på att metoden anrop kör .NET **CallMethodOnDevice** app att anropa metoden i appen simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="11b3e-153">Now that the device is connected and waiting for method invocations, run the .NET **CallMethodOnDevice** app to invoke the method in the simulated device app.</span></span> <span data-ttu-id="11b3e-154">Du bör se svaret från enheten som har skrivits i konsolen.</span><span class="sxs-lookup"><span data-stu-id="11b3e-154">You should see the device response written in the console.</span></span>
   
    ![][8]
4. <span data-ttu-id="11b3e-155">Enheten reagerar sedan till metoden genom att skriva ut det här meddelandet:</span><span class="sxs-lookup"><span data-stu-id="11b3e-155">The device then reacts to the method by printing this message:</span></span>
   
    ![][9]

## <a name="next-steps"></a><span data-ttu-id="11b3e-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="11b3e-156">Next steps</span></span>
<span data-ttu-id="11b3e-157">I den här självstudiekursen konfigurerade du en ny IoT Hub på Azure Portal och skapade sedan en enhetsidentitet i IoT-hubbens identitetsregister.</span><span class="sxs-lookup"><span data-stu-id="11b3e-157">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="11b3e-158">Denna enhetsidentitet använde för att aktivera appen simulerade enheten att ta hänsyn till metoder som anropas av molnet.</span><span class="sxs-lookup"><span data-stu-id="11b3e-158">You used this device identity to enable the simulated device app to react to methods invoked by the cloud.</span></span> <span data-ttu-id="11b3e-159">Du kan också skapat en app som anropar metoder på enheten och visar svaret från enheten.</span><span class="sxs-lookup"><span data-stu-id="11b3e-159">You also created an app that invokes methods on the device and displays the response from the device.</span></span> 

<span data-ttu-id="11b3e-160">Mer information om hur du kan komma igång med IoT Hub och utforska andra IoT-scenarier finns här:</span><span class="sxs-lookup"><span data-stu-id="11b3e-160">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="11b3e-161">[Kom igång med IoT-hubb]</span><span class="sxs-lookup"><span data-stu-id="11b3e-161">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="11b3e-162">[Schema-jobb på flera enheter][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="11b3e-162">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="11b3e-163">Information om hur du utökar din IoT-lösningen och schema metodanrop på flera enheter finns i [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="11b3e-163">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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
<span data-ttu-id="11b3e-164">[Kom igång med IoT-hubb]: iot-hub-node-node-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="11b3e-164">[Get started with IoT Hub]: iot-hub-node-node-getstarted.md</span></span>
