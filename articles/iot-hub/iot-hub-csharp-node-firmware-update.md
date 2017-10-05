---
title: Enheten firmware-uppdatering med Azure IoT Hub (.NET/nod) | Microsoft Docs
description: "Hur du använder hantering av enheter på Azure IoT Hub för att initiera en firmware-uppdatering för enheten. Du kan använda Azure IoT-enhet SDK för Node.js för att implementera en simulerad enhetsapp och tjänsten Azure IoT SDK för .NET att implementera ett service-appen som utlöser firmware-uppdatering."
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
ms.openlocfilehash: c2192328a152e955d182c4a07b391c98a5960964
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnode"></a><span data-ttu-id="f9b02-104">Använd enhetshantering för att starta en enhet på en firmware-uppdatering (.NET/nod)</span><span class="sxs-lookup"><span data-stu-id="f9b02-104">Use device management to initiate a device firmware update (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a><span data-ttu-id="f9b02-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="f9b02-105">Introduction</span></span>
<span data-ttu-id="f9b02-106">I den [Kom igång med enhetshantering] [ lnk-dm-getstarted] självstudiekursen du sett hur du använder den [enheten dubbla] [ lnk-devtwin] och [direkt metoder] [ lnk-c2dmethod] primitiver att starta om en enhet via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="f9b02-106">In the [Get started with device management][lnk-dm-getstarted] tutorial, you saw how to use the [device twin][lnk-devtwin] and [direct methods][lnk-c2dmethod] primitives to remotely reboot a device.</span></span> <span data-ttu-id="f9b02-107">Den här kursen visar hur du gör en slutpunkt till slutpunkt simulerade firmware-uppdatering använder samma IoT-hubb primitiver.</span><span class="sxs-lookup"><span data-stu-id="f9b02-107">This tutorial uses the same IoT Hub primitives and shows you how to do an end-to-end simulated firmware update.</span></span>  <span data-ttu-id="f9b02-108">Det här mönstret används i uppdatering av inbyggd hanteringsprogramvara för den [hallon Pi enheten implementering exempel][lnk-rpi-implementation].</span><span class="sxs-lookup"><span data-stu-id="f9b02-108">This pattern is used in the firmware update implementation for the [Raspberry Pi device implementation sample][lnk-rpi-implementation].</span></span>

<span data-ttu-id="f9b02-109">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="f9b02-109">This tutorial shows you how to:</span></span>

* <span data-ttu-id="f9b02-110">Skapa en .NET-konsolapp som anropar metoden firmwareUpdate direkt i appen simulerade enheten via din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f9b02-110">Create a .NET console app that calls the firmwareUpdate direct method in the simulated device app through your IoT hub.</span></span>
* <span data-ttu-id="f9b02-111">Skapa en simulerad enhetsapp som implementerar en **firmwareUpdate** direkt metod.</span><span class="sxs-lookup"><span data-stu-id="f9b02-111">Create a simulated device app that implements a **firmwareUpdate** direct method.</span></span> <span data-ttu-id="f9b02-112">Den här metoden initierar en process i flera steg som väntar på att ladda ned avbildningen inbyggd programvara, laddar ned avbildningen av inbyggd programvara och slutligen använder inbyggd avbildningen.</span><span class="sxs-lookup"><span data-stu-id="f9b02-112">This method initiates a multi-stage process that waits to download the firmware image, downloads the firmware image, and finally applies the firmware image.</span></span> <span data-ttu-id="f9b02-113">Under varje steg i uppdateringen använder enheten rapporterade egenskaperna för att rapportera förlopp.</span><span class="sxs-lookup"><span data-stu-id="f9b02-113">During each stage of the update, the device uses the reported properties to report on progress.</span></span>

<span data-ttu-id="f9b02-114">I slutet av den här kursen har du en enhet för Node.js konsolapp och en serverdel för .NET (C#) konsolapp:</span><span class="sxs-lookup"><span data-stu-id="f9b02-114">At the end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="f9b02-115">**dmpatterns_fwupdate_service.js**, som anropar en direkt metod i appen simulerade enheten visar svaret och regelbundet (varje 500ms) visar den uppdaterade rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f9b02-115">**dmpatterns_fwupdate_service.js**, which calls a direct method in the simulated device app, displays the response, and periodically (every 500ms) displays the updated reported properties.</span></span>

<span data-ttu-id="f9b02-116">**TriggerFWUpdate**, som ansluter till din IoT-hubb med enhetens identitet skapade tidigare, tar emot en firmwareUpdate direkt metod, körs via en process som flera tillstånd för att simulera en firmware-uppdatering inklusive: väntar på avbildningen hämtas, hämta den nya avbildningen och slutligen avbildningen används.</span><span class="sxs-lookup"><span data-stu-id="f9b02-116">**TriggerFWUpdate**, which connects to your IoT hub with the device identity created earlier, receives a firmwareUpdate direct method, runs through a multi-state process to simulate a firmware update including: waiting for the image download, downloading the new image, and finally applying the image.</span></span>

<span data-ttu-id="f9b02-117">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f9b02-117">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="f9b02-118">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="f9b02-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="f9b02-119">Node.js-version 0.12.x eller senare</span><span class="sxs-lookup"><span data-stu-id="f9b02-119">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="f9b02-120">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur du installerar Node.js för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="f9b02-120">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="f9b02-121">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="f9b02-121">An active Azure account.</span></span> <span data-ttu-id="f9b02-122">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="f9b02-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="f9b02-123">Följ den [Kom igång med enhetshantering](iot-hub-csharp-node-device-management-get-started.md) artikel för att skapa din IoT-hubb och hämta anslutningssträngen för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f9b02-123">Follow the [Get started with device management](iot-hub-csharp-node-device-management-get-started.md) article to create your IoT hub and get your IoT Hub connection string.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a><span data-ttu-id="f9b02-124">Utlös en fjärransluten firmware-uppdatering på enheten med en direkt metod</span><span class="sxs-lookup"><span data-stu-id="f9b02-124">Trigger a remote firmware update on the device using a direct method</span></span>
<span data-ttu-id="f9b02-125">I det här avsnittet skapar du en .NET-konsolapp (med C#) som initierar en fjärransluten firmware-uppdatering på en enhet.</span><span class="sxs-lookup"><span data-stu-id="f9b02-125">In this section, you create a .NET console app (using C#) that initiates a remote firmware update on a device.</span></span> <span data-ttu-id="f9b02-126">Appen använder direkta metoden för att initiera uppdateringen och använder enheten dubbla frågor för att hämta status för aktiva firmware-uppdatering med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="f9b02-126">The app uses a direct method to initiate the update and uses device twin queries to periodically get the status of the active firmware update.</span></span>

1. <span data-ttu-id="f9b02-127">I Visual Studio lägger du till ett Visual C# Classic Desktop-projekt i den aktuella lösningen med hjälp av projektmallen **Konsolprogram**.</span><span class="sxs-lookup"><span data-stu-id="f9b02-127">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="f9b02-128">Namnge projektet **TriggerFWUpdate**.</span><span class="sxs-lookup"><span data-stu-id="f9b02-128">Name the project **TriggerFWUpdate**.</span></span>

    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]

1. <span data-ttu-id="f9b02-130">I Solution Explorer högerklickar du på den **TriggerFWUpdate** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="f9b02-130">In Solution Explorer, right-click the **TriggerFWUpdate** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="f9b02-131">Välj **Bläddra** i fönstret för **NuGet-pakethanteraren**, leta upp **microsoft.azure.devices**, välj **Installera** för att installera **Microsoft.Azure.Devices**-paketet och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="f9b02-131">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="f9b02-132">Denna procedur hämtar, installerar och lägger till en referens för [NuGet-paketetet SDK för Azure IoT-tjänster][lnk-nuget-service-sdk] och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="f9b02-132">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. <span data-ttu-id="f9b02-134">Lägg till följande `using`-uttryck överst i **Program.cs**-filen:</span><span class="sxs-lookup"><span data-stu-id="f9b02-134">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. <span data-ttu-id="f9b02-135">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="f9b02-135">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="f9b02-136">Ersätt flera platshållarvärdena med IoT-hubb anslutningssträngen för hubben som du skapade i föregående avsnitt och Id för din enhet.</span><span class="sxs-lookup"><span data-stu-id="f9b02-136">Replace the multiple placeholder values with the IoT Hub connection string for the hub that you created in the previous section and the Id of your device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. <span data-ttu-id="f9b02-137">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="f9b02-137">Add the following method to the **Program** class:</span></span>
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. <span data-ttu-id="f9b02-138">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="f9b02-138">Add the following method to the **Program** class:</span></span>

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

1. <span data-ttu-id="f9b02-139">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="f9b02-139">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
1. <span data-ttu-id="f9b02-140">I Solution Explorer, öppna den **ange Startprojekt...**  och kontrollera att den **åtgärd** för **TriggerFWUpdate** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="f9b02-140">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **TriggerFWUpdate** project is **Start**.</span></span>

1. <span data-ttu-id="f9b02-141">Skapa lösningen.</span><span class="sxs-lookup"><span data-stu-id="f9b02-141">Build the solution.</span></span>

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a><span data-ttu-id="f9b02-142">Kör apparna</span><span class="sxs-lookup"><span data-stu-id="f9b02-142">Run the apps</span></span>
<span data-ttu-id="f9b02-143">Nu är det dags att köra apparna.</span><span class="sxs-lookup"><span data-stu-id="f9b02-143">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="f9b02-144">I Kommandotolken i den **manageddevice** mapp, kör följande kommando för att börja lyssna efter omstart direkta metoden.</span><span class="sxs-lookup"><span data-stu-id="f9b02-144">At the command prompt in the **manageddevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. <span data-ttu-id="f9b02-145">I Visual Studio högerklickar du på den **TriggerFWUpdate** projectRun till C#-konsolen appen, Välj **felsöka** och **Starta ny instans**.</span><span class="sxs-lookup"><span data-stu-id="f9b02-145">In Visual Studio, right-click on the **TriggerFWUpdate** projectRun to the C# console app, select **Debug** and **Start new instance**.</span></span>

3. <span data-ttu-id="f9b02-146">Svaret från enheten till den direkta metoden i konsolen visas.</span><span class="sxs-lookup"><span data-stu-id="f9b02-146">You see the device response to the direct method in the console.</span></span>

    ![Inbyggd programvara har uppdaterats][img-fwupdate]

## <a name="next-steps"></a><span data-ttu-id="f9b02-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f9b02-148">Next steps</span></span>
<span data-ttu-id="f9b02-149">I den här kursen används direkt metod för att utlösa en fjärransluten firmware-uppdatering på en enhet och används egenskaperna rapporterade för att följa förloppet för firmware-uppdatering.</span><span class="sxs-lookup"><span data-stu-id="f9b02-149">In this tutorial, you used a direct method to trigger a remote firmware update on a device and used the reported properties to follow the progress of the firmware update.</span></span>

<span data-ttu-id="f9b02-150">Information om hur du utökar din IoT-lösningen och schema metodanrop på flera enheter finns i [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="f9b02-150">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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