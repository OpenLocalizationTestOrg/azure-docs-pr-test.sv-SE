---
title: aaaDevice firmware-uppdatering med Azure IoT Hub (.NET/nod) | Microsoft Docs
description: "Hur uppdaterar toouse enhetshantering i Azure IoT Hub tooinitiate en enhetens inbyggda programvara. Du använder hello Azure IoT-enhet SDK för Node.js tooimplement en simulerad enhetsapp och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som utlöser hello firmware-uppdatering."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/17/2017
ms.author: juanpere
ms.openlocfilehash: d48899651bcd5d67e577c971be0f9453c8f21592
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-device-management-tooinitiate-a-device-firmware-update-netnode"></a><span data-ttu-id="83ee0-104">Använda device management tooinitiate en enhet på en firmware-uppdatering (.NET/nod)</span><span class="sxs-lookup"><span data-stu-id="83ee0-104">Use device management tooinitiate a device firmware update (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a><span data-ttu-id="83ee0-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="83ee0-105">Introduction</span></span>
<span data-ttu-id="83ee0-106">I hello [komma igång med hantering av enheter] [ lnk-dm-getstarted] självstudiekursen kommer du sett hur toouse hello [enheten dubbla] [ lnk-devtwin] och [direkt metoder] [ lnk-c2dmethod] primitiver tooremotely starta om en enhet.</span><span class="sxs-lookup"><span data-stu-id="83ee0-106">In hello [Get started with device management][lnk-dm-getstarted] tutorial, you saw how toouse hello [device twin][lnk-devtwin] and [direct methods][lnk-c2dmethod] primitives tooremotely reboot a device.</span></span> <span data-ttu-id="83ee0-107">Den här självstudiekursen använder hello samma IoT-hubb primitiver och visar hur toodo en slutpunkt till slutpunkt simulerade firmware-uppdatering.</span><span class="sxs-lookup"><span data-stu-id="83ee0-107">This tutorial uses hello same IoT Hub primitives and shows you how toodo an end-to-end simulated firmware update.</span></span>  <span data-ttu-id="83ee0-108">Det här mönstret används i hello uppdatering av inbyggd hanteringsprogramvara för hello [hallon Pi enheten implementering exempel][lnk-rpi-implementation].</span><span class="sxs-lookup"><span data-stu-id="83ee0-108">This pattern is used in hello firmware update implementation for hello [Raspberry Pi device implementation sample][lnk-rpi-implementation].</span></span>

<span data-ttu-id="83ee0-109">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="83ee0-109">This tutorial shows you how to:</span></span>

* <span data-ttu-id="83ee0-110">Skapa en .NET-konsolapp som anropar hello firmwareUpdate direkt metod i hello simulerade enheten appen via din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="83ee0-110">Create a .NET console app that calls hello firmwareUpdate direct method in hello simulated device app through your IoT hub.</span></span>
* <span data-ttu-id="83ee0-111">Skapa en simulerad enhetsapp som implementerar en **firmwareUpdate** direkt metod.</span><span class="sxs-lookup"><span data-stu-id="83ee0-111">Create a simulated device app that implements a **firmwareUpdate** direct method.</span></span> <span data-ttu-id="83ee0-112">Den här metoden initierar en process i flera steg som väntar toodownload hello firmware bild, laddar hello firmware avbildningen och slutligen gäller hello firmware avbildningen.</span><span class="sxs-lookup"><span data-stu-id="83ee0-112">This method initiates a multi-stage process that waits toodownload hello firmware image, downloads hello firmware image, and finally applies hello firmware image.</span></span> <span data-ttu-id="83ee0-113">Under varje steg i hello update rapporterade hello enheten använder hello egenskaper tooreport på pågår.</span><span class="sxs-lookup"><span data-stu-id="83ee0-113">During each stage of hello update, hello device uses hello reported properties tooreport on progress.</span></span>

<span data-ttu-id="83ee0-114">Hello slutet av den här självstudiekursen har du en enhet för Node.js konsolapp och en serverdel för .NET (C#) konsolapp:</span><span class="sxs-lookup"><span data-stu-id="83ee0-114">At hello end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="83ee0-115">**dmpatterns_fwupdate_service.js**, som anropar en direkt metod i hello simulerade enhetsapp visar hello svar och regelbundet (varje 500ms) visar hello uppdateras rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="83ee0-115">**dmpatterns_fwupdate_service.js**, which calls a direct method in hello simulated device app, displays hello response, and periodically (every 500ms) displays hello updated reported properties.</span></span>

<span data-ttu-id="83ee0-116">**TriggerFWUpdate**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare, tar emot en firmwareUpdate direkt metod, körs via en flera tillstånd processen toosimulate inklusive en inbyggd uppdatering: väntar på hello avbildningen hämtas hämtar hello ny avbildning och slutligen använder hello-bild.</span><span class="sxs-lookup"><span data-stu-id="83ee0-116">**TriggerFWUpdate**, which connects tooyour IoT hub with hello device identity created earlier, receives a firmwareUpdate direct method, runs through a multi-state process toosimulate a firmware update including: waiting for hello image download, downloading hello new image, and finally applying hello image.</span></span>

<span data-ttu-id="83ee0-117">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="83ee0-117">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="83ee0-118">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="83ee0-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="83ee0-119">Node.js-version 0.12.x eller senare</span><span class="sxs-lookup"><span data-stu-id="83ee0-119">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="83ee0-120">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="83ee0-120">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="83ee0-121">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="83ee0-121">An active Azure account.</span></span> <span data-ttu-id="83ee0-122">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="83ee0-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="83ee0-123">Följ hello [Kom igång med enhetshantering](iot-hub-csharp-node-device-management-get-started.md) artikel toocreate din IoT-hubb och hämta anslutningssträngen för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="83ee0-123">Follow hello [Get started with device management](iot-hub-csharp-node-device-management-get-started.md) article toocreate your IoT hub and get your IoT Hub connection string.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a><span data-ttu-id="83ee0-124">Utlös en fjärransluten firmware-uppdatering på hello-enhet med en direkt metod</span><span class="sxs-lookup"><span data-stu-id="83ee0-124">Trigger a remote firmware update on hello device using a direct method</span></span>
<span data-ttu-id="83ee0-125">I det här avsnittet skapar du en .NET-konsolapp (med C#) som initierar en fjärransluten firmware-uppdatering på en enhet.</span><span class="sxs-lookup"><span data-stu-id="83ee0-125">In this section, you create a .NET console app (using C#) that initiates a remote firmware update on a device.</span></span> <span data-ttu-id="83ee0-126">hello appen använder en direkta metoden tooinitiate hello uppdatering och använder enheten dubbla frågor tooperiodically få hello status för hello active firmware-uppdatering.</span><span class="sxs-lookup"><span data-stu-id="83ee0-126">hello app uses a direct method tooinitiate hello update and uses device twin queries tooperiodically get hello status of hello active firmware update.</span></span>

1. <span data-ttu-id="83ee0-127">I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="83ee0-127">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="83ee0-128">Namnet hello projektet **TriggerFWUpdate**.</span><span class="sxs-lookup"><span data-stu-id="83ee0-128">Name hello project **TriggerFWUpdate**.</span></span>

    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]

1. <span data-ttu-id="83ee0-130">I Solution Explorer högerklickar du på hello **TriggerFWUpdate** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="83ee0-130">In Solution Explorer, right-click hello **TriggerFWUpdate** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="83ee0-131">I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="83ee0-131">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="83ee0-132">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="83ee0-132">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. <span data-ttu-id="83ee0-134">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="83ee0-134">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. <span data-ttu-id="83ee0-135">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="83ee0-135">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="83ee0-136">Ersätt hello flera platshållarvärdena med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello och hello-Id för din enhet.</span><span class="sxs-lookup"><span data-stu-id="83ee0-136">Replace hello multiple placeholder values with hello IoT Hub connection string for hello hub that you created in hello previous section and hello Id of your device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. <span data-ttu-id="83ee0-137">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="83ee0-137">Add hello following method toohello **Program** class:</span></span>
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. <span data-ttu-id="83ee0-138">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="83ee0-138">Add hello following method toohello **Program** class:</span></span>

        public static async Task StartFirmwareUpdate()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);
            method.SetPayloadJson(
                @"{
                    fwPackageUri : 'https://someurl'
                }");

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

1. <span data-ttu-id="83ee0-139">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="83ee0-139">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
1. <span data-ttu-id="83ee0-140">Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **TriggerFWUpdate** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="83ee0-140">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **TriggerFWUpdate** project is **Start**.</span></span>

1. <span data-ttu-id="83ee0-141">Skapa hello lösning.</span><span class="sxs-lookup"><span data-stu-id="83ee0-141">Build hello solution.</span></span>

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a><span data-ttu-id="83ee0-142">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="83ee0-142">Run hello apps</span></span>
<span data-ttu-id="83ee0-143">Du är nu redo toorun hello appar.</span><span class="sxs-lookup"><span data-stu-id="83ee0-143">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="83ee0-144">Kommandotolken hello i hello **manageddevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.</span><span class="sxs-lookup"><span data-stu-id="83ee0-144">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. <span data-ttu-id="83ee0-145">I Visual Studio högerklickar du på hello **TriggerFWUpdate** projectRun toohello C#-konsolapp, Välj **felsöka** och **Starta ny instans**.</span><span class="sxs-lookup"><span data-stu-id="83ee0-145">In Visual Studio, right-click on hello **TriggerFWUpdate** projectRun toohello C# console app, select **Debug** and **Start new instance**.</span></span>

3. <span data-ttu-id="83ee0-146">Du ser hello enheten svar toohello direkt metod i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="83ee0-146">You see hello device response toohello direct method in hello console.</span></span>

    ![Inbyggd programvara har uppdaterats][img-fwupdate]

## <a name="next-steps"></a><span data-ttu-id="83ee0-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="83ee0-148">Next steps</span></span>
<span data-ttu-id="83ee0-149">I kursen får du använde en direkta metoden tootrigger en fjärransluten firmware-uppdatering på en enhet och använda hello rapporterade egenskaper toofollow hello fortskrider hello firmware-uppdatering.</span><span class="sxs-lookup"><span data-stu-id="83ee0-149">In this tutorial, you used a direct method tootrigger a remote firmware update on a device and used hello reported properties toofollow hello progress of hello firmware update.</span></span>

<span data-ttu-id="83ee0-150">toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="83ee0-150">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-firmware-update/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-firmware-update/createnetapp.png
[img-fwupdate]: media/iot-hub-csharp-node-firmware-update/fwupdated.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/