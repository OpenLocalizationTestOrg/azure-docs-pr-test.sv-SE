---
title: aaaUse Azure IoT Hub direkt metoder (.NET/.NET) | Microsoft Docs
description: "Hur toouse Azure IoT Hub direkt metoder. Du kan använda hello Azure IoT-enhet SDK för .NET tooimplement en simulerad enhetsapp som innehåller en direkt metod och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som anropar hello direkta metoden."
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: dkshir
ms.openlocfilehash: d4fa093a99558ec6faf294c2583a14a722b9ac03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-netnet"></a><span data-ttu-id="2b549-104">Använda direkt metoder (.NET/.NET)</span><span class="sxs-lookup"><span data-stu-id="2b549-104">Use direct methods (.NET/.NET)</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="2b549-105">I den här kursen är vi kommer toodevelop två .NET konsolappar:</span><span class="sxs-lookup"><span data-stu-id="2b549-105">In this tutorial, we are going toodevelop two .NET console apps:</span></span>

* <span data-ttu-id="2b549-106">**CallMethodOnDevice**, en backend-app, som anropar en metod i hello simulerade enhetsapp och visar hello svar.</span><span class="sxs-lookup"><span data-stu-id="2b549-106">**CallMethodOnDevice**, a back-end app, which calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="2b549-107">**SimulateDeviceMethods**, en konsolapp som simulerar en enhet som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och svarar toohello-metoden anropas av hello molnet.</span><span class="sxs-lookup"><span data-stu-id="2b549-107">**SimulateDeviceMethods**, a console app which simulates a device connecting tooyour IoT hub with hello device identity created earlier, and responds toohello method called by hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="2b549-108">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild toorun både program på enheter och din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="2b549-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="2b549-109">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="2b549-109">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="2b549-110">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2b549-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="2b549-111">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="2b549-111">An active Azure account.</span></span> <span data-ttu-id="2b549-112">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="2b549-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="2b549-113">Om du vill toocreate hello enhetsidentitet programmässigt i stället kan du läsa hello motsvarande avsnitt i hello [ansluta din simulerade enhet tooyour IoT-hubb med hjälp av .NET] [ lnk-device-identity-csharp] artikel.</span><span class="sxs-lookup"><span data-stu-id="2b549-113">If you want toocreate hello device identity programmatically instead, read hello corresponding section in hello [Connect your simulated device tooyour IoT hub using .NET][lnk-device-identity-csharp] article.</span></span>


## <a name="create-a-simulated-device-app"></a><span data-ttu-id="2b549-114">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="2b549-114">Create a simulated device app</span></span>
<span data-ttu-id="2b549-115">I det här avsnittet skapar du en .NET-konsolapp som svarar tooa metoden anropas av hello-lösningen slutet.</span><span class="sxs-lookup"><span data-stu-id="2b549-115">In this section, you create a .NET console app that responds tooa method called by hello solution back end.</span></span>

1. <span data-ttu-id="2b549-116">I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="2b549-116">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="2b549-117">Namnet hello projektet **SimulateDeviceMethods**.</span><span class="sxs-lookup"><span data-stu-id="2b549-117">Name hello project **SimulateDeviceMethods**.</span></span>
   
    ![Ny Visual C# Windows Classic enhetsapp][img-createdeviceapp]
    
1. <span data-ttu-id="2b549-119">I Solution Explorer högerklickar du på hello **SimulateDeviceMethods** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="2b549-119">In Solution Explorer, right-click hello **SimulateDeviceMethods** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="2b549-120">I hello **NuGet Package Manager** väljer **Bläddra** och Sök efter **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="2b549-120">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="2b549-121">Välj **installera** tooinstall hello **Microsoft.Azure.Devices.Client** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="2b549-121">Select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="2b549-122">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT-enhet SDK] [ lnk-nuget-client-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="2b549-122">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet-Pakethanteraren fönstret-klientappen][img-clientnuget]
1. <span data-ttu-id="2b549-124">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="2b549-124">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;

1. <span data-ttu-id="2b549-125">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="2b549-125">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="2b549-126">Ersätt hello platshållarvärde med anslutningssträngen för hello enheten som du antecknade i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2b549-126">Replace hello placeholder value with hello device connection string that you noted in hello previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. <span data-ttu-id="2b549-127">Lägg till hello följande tooimplement hello direkt metod på hello enhet:</span><span class="sxs-lookup"><span data-stu-id="2b549-127">Add hello following tooimplement hello direct method on hello device:</span></span>

        static Task<MethodResponse> WriteLineToConsole(MethodRequest methodRequest, object userContext)
        {
            Console.WriteLine();
            Console.WriteLine("\t{0}", methodRequest.DataAsJson);
            Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);

            string result = "'Input was written toolog.'";
            return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
        }

1. <span data-ttu-id="2b549-128">Slutligen lägger du till följande kod toohello hello **Main** metoden tooopen hello anslutning tooyour IoT hub och initiera hello metoden lyssnare:</span><span class="sxs-lookup"><span data-stu-id="2b549-128">Finally, add hello following code toohello **Main** method tooopen hello connection tooyour IoT hub and initialize hello method listener:</span></span>
   
        try
        {
            Console.WriteLine("Connecting toohub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

            // setup callback for "writeLine" method
            Client.SetMethodHandlerAsync("writeLine", WriteLineToConsole, null).Wait();
            Console.WriteLine("Waiting for direct method call\n Press enter tooexit.");
            Console.ReadLine();

            Console.WriteLine("Exiting...");

            // as a good practice, remove hello "writeLine" handler
            Client.SetMethodHandlerAsync("writeLine", null, null).Wait();
            Client.CloseAsync().Wait();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
        
1. <span data-ttu-id="2b549-129">I hello Visual Studio Solution Explorer högerklickar du på din lösning och klicka sedan på **ange Startprojekt...** . Välj **enda Startprojekt**, och välj sedan hello **SimulateDeviceMethods** projekt i hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="2b549-129">In hello Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**. Select **Single startup project**, and then select hello **SimulateDeviceMethods** project in hello dropdown menu.</span></span>        

> [!NOTE]
> <span data-ttu-id="2b549-130">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="2b549-130">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="2b549-131">I produktionskod, bör du implementera försök principer (till exempel anslutning försök), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="2b549-131">In production code, you should implement retry policies (such as connection retry), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="2b549-132">Anropa en metod som direkt på en enhet</span><span class="sxs-lookup"><span data-stu-id="2b549-132">Call a direct method on a device</span></span>
<span data-ttu-id="2b549-133">I det här avsnittet skapar du en .NET-konsolapp som anropar en metod i hello simulerade enheten appen och sedan visar hello svar.</span><span class="sxs-lookup"><span data-stu-id="2b549-133">In this section, you create a .NET console app that calls a method in hello simulated device app and then displays hello response.</span></span>

1. <span data-ttu-id="2b549-134">I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="2b549-134">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="2b549-135">Se till att hello av .NET Framework 4.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2b549-135">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="2b549-136">Namnet hello projektet **CallMethodOnDevice**.</span><span class="sxs-lookup"><span data-stu-id="2b549-136">Name hello project **CallMethodOnDevice**.</span></span>
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createserviceapp]
2. <span data-ttu-id="2b549-138">I Solution Explorer högerklickar du på hello **CallMethodOnDevice** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="2b549-138">In Solution Explorer, right-click hello **CallMethodOnDevice** project, and then click **Manage NuGet Packages...**.</span></span>
3. <span data-ttu-id="2b549-139">I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="2b549-139">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="2b549-140">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="2b549-140">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]

4. <span data-ttu-id="2b549-142">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="2b549-142">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="2b549-143">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="2b549-143">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="2b549-144">Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="2b549-144">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="2b549-145">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="2b549-145">Add hello following method toohello **Program** class:</span></span>
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line toobe written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    <span data-ttu-id="2b549-146">Den här metoden startar direkt metod med namnet `writeLine` på hello `myDeviceId` enhet.</span><span class="sxs-lookup"><span data-stu-id="2b549-146">This method invokes a direct method with name `writeLine` on hello `myDeviceId` device.</span></span> <span data-ttu-id="2b549-147">Sedan skriver hello-svar som tillhandahålls av hello enheten på hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="2b549-147">Then, it writes hello response provided by hello device on hello console.</span></span> <span data-ttu-id="2b549-148">Observera hur det är möjligt toospecify ett timeout-värde för hello enheten toorespond.</span><span class="sxs-lookup"><span data-stu-id="2b549-148">Note how it is possible toospecify a timeout value for hello device toorespond.</span></span>
7. <span data-ttu-id="2b549-149">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="2b549-149">Finally, add hello following lines toohello **Main** method:</span></span>
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. <span data-ttu-id="2b549-150">I hello Visual Studio Solution Explorer högerklickar du på din lösning och klicka sedan på **ange Startprojekt...** . Välj **enda Startprojekt**, och välj sedan hello **CallMethodOnDevice** projekt i hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="2b549-150">In hello Visual Studio Solution Explorer, right-click your solution, and then click **Set StartUp Projects...**. Select **Single startup project**, and then select hello **CallMethodOnDevice** project in hello dropdown menu.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="2b549-151">Köra hello program</span><span class="sxs-lookup"><span data-stu-id="2b549-151">Run hello applications</span></span>
<span data-ttu-id="2b549-152">Du är nu redo toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="2b549-152">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="2b549-153">Kör hello .NET enhetsapp **SimulateDeviceMethods**.</span><span class="sxs-lookup"><span data-stu-id="2b549-153">Run hello .NET device app **SimulateDeviceMethods**.</span></span> <span data-ttu-id="2b549-154">Den ska börja lyssna efter metodanrop från din IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="2b549-154">It should start listening for method calls from your IoT Hub:</span></span> 

    ![Enhetsapp kör][img-deviceapprun]
1. <span data-ttu-id="2b549-156">Nu hello enheten är ansluten och väntar på att metoden anrop, kör hello .NET **CallMethodOnDevice** tooinvoke hello mobilappmetoden i hello simulerade enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="2b549-156">Now that hello device is connected and waiting for method invocations, run hello .NET **CallMethodOnDevice** app tooinvoke hello method in hello simulated device app.</span></span> <span data-ttu-id="2b549-157">Du bör se hello enhetens svar skrivs i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="2b549-157">You should see hello device response written in hello console.</span></span>
   
    ![Kör Service-appen][img-serviceapprun]
1. <span data-ttu-id="2b549-159">hello enheten reagerar sedan toohello metoden genom att skriva ut det här meddelandet:</span><span class="sxs-lookup"><span data-stu-id="2b549-159">hello device then reacts toohello method by printing this message:</span></span>
   
    ![Direkta metoden anropas på hello-enhet][img-directmethodinvoked]

## <a name="next-steps"></a><span data-ttu-id="2b549-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b549-161">Next steps</span></span>
<span data-ttu-id="2b549-162">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="2b549-162">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="2b549-163">Du har använt den här enheten identitet tooenable hello simulerade enheten app tooreact toomethods anropas av hello molnet.</span><span class="sxs-lookup"><span data-stu-id="2b549-163">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="2b549-164">Skapade du även en app som anropar metoder på hello enhet och visar hello svar från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="2b549-164">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span> 

<span data-ttu-id="2b549-165">toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:</span><span class="sxs-lookup"><span data-stu-id="2b549-165">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="2b549-166">[Kom igång med IoT-hubb]</span><span class="sxs-lookup"><span data-stu-id="2b549-166">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="2b549-167">[Schema-jobb på flera enheter][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="2b549-167">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="2b549-168">toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="2b549-168">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

<!-- Images. -->
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-device-app.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-direct-methods/device-app-nuget.png
[img-createserviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-service-app.png
[img-servicenuget]: ./media/iot-hub-csharp-csharp-direct-methods/service-app-nuget.png
[img-deviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-device-app.png
[img-serviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-service-app.png
[img-directmethodinvoked]: ./media/iot-hub-csharp-csharp-direct-methods/direct-method-invoked.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[Kom igång med IoT-hubb]: iot-hub-node-node-getstarted.md
