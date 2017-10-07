---
title: "aaaGet igång med Azure IoT Hub-enhet twins (.NET/nod) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet twins tooadd taggar och sedan använda en IoT-hubb-fråga. Du kan använda hello Azure IoT-enhet SDK för Node.js tooimplement hello simulerade enheten appen och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som lägger till hello taggar och kör hello IoT-hubb frågan."
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: 
ms.assetid: f7e23b6e-bfde-4fba-a6ec-dbb0f0e005f4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: elioda
ms.openlocfilehash: 1cec082ebddc19c9b87998a5fd0159d32b07acd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-netnode"></a><span data-ttu-id="bd687-104">Kom igång med enheten twins (.NET/nod)</span><span class="sxs-lookup"><span data-stu-id="bd687-104">Get started with device twins (.NET/Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="bd687-105">Hello slutet av den här självstudiekursen har du en .NET och Node.js-konsolapp:</span><span class="sxs-lookup"><span data-stu-id="bd687-105">At hello end of this tutorial, you will have a .NET and a Node.js console app:</span></span>

* <span data-ttu-id="bd687-106">**AddTagsAndQuery.sln**, en .NET backend-app som lägger till taggar och frågar enheten twins.</span><span class="sxs-lookup"><span data-stu-id="bd687-106">**AddTagsAndQuery.sln**, a .NET back-end app, which adds tags and queries device twins.</span></span>
* <span data-ttu-id="bd687-107">**TwinSimulatedDevice.js**, en Node.js-app som simulerar en enhet som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och rapporterar tillståndet anslutningen.</span><span class="sxs-lookup"><span data-stu-id="bd687-107">**TwinSimulatedDevice.js**, a Node.js app which simulates a device that connects tooyour IoT hub with hello device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="bd687-108">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="bd687-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="bd687-109">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="bd687-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="bd687-110">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="bd687-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="bd687-111">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="bd687-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="bd687-112">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="bd687-112">An active Azure account.</span></span> <span data-ttu-id="bd687-113">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="bd687-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-service-app"></a><span data-ttu-id="bd687-114">Skapa hello service-appen</span><span class="sxs-lookup"><span data-stu-id="bd687-114">Create hello service app</span></span>
<span data-ttu-id="bd687-115">I det här avsnittet skapar du en .NET-konsolapp (med C#) som lägger till platsen metadata toohello enheten dubbla som är associerade med **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="bd687-115">In this section, you create a .NET console app (using C#) that adds location metadata toohello device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="bd687-116">Därefter frågor hello enheten twins lagras i hello IoT-hubb hello enheter finns i hello oss och hello som rapporterats av en mobil anslutning.</span><span class="sxs-lookup"><span data-stu-id="bd687-116">It then queries hello device twins stored in hello IoT hub selecting hello devices located in hello US, and then hello ones that reported a cellular connection.</span></span>

1. <span data-ttu-id="bd687-117">I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="bd687-117">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="bd687-118">Namnet hello projektet **AddTagsAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="bd687-118">Name hello project **AddTagsAndQuery**.</span></span>
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]
1. <span data-ttu-id="bd687-120">I Solution Explorer högerklickar du på hello **AddTagsAndQuery** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="bd687-120">In Solution Explorer, right-click hello **AddTagsAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="bd687-121">I hello **NuGet Package Manager** väljer **Bläddra** och Sök efter **microsoft.azure.devices**.</span><span class="sxs-lookup"><span data-stu-id="bd687-121">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices**.</span></span> <span data-ttu-id="bd687-122">Välj **installera** tooinstall hello **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="bd687-122">Select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="bd687-123">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="bd687-123">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. <span data-ttu-id="bd687-125">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="bd687-125">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
1. <span data-ttu-id="bd687-126">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="bd687-126">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="bd687-127">Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="bd687-127">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="bd687-128">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="bd687-128">Add hello following method toohello **Program** class:</span></span>
   
        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }
   
    <span data-ttu-id="bd687-129">Hej **RegistryManager** klassen visar alla hello metoder krävs toointeract med enheten twins från hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bd687-129">hello **RegistryManager** class exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="bd687-130">hello föregående kod först initierar hello **registryManager** objekt, och sedan hämtar hello enheten dubbla för **myDeviceId**, och slutligen uppdateras taggarna hello önskad platsinformation.</span><span class="sxs-lookup"><span data-stu-id="bd687-130">hello previous code first initializes hello **registryManager** object, then retrieves hello device twin for **myDeviceId**, and finally updates its tags with hello desired location information.</span></span>
   
    <span data-ttu-id="bd687-131">När du har uppdaterat, körs två frågor: hello väljer först endast hello enheten twins av enheter som finns i hello **Redmond43** anläggningar och hello andra förfinar hello frågan tooselect bara hello enheter som även är anslutna via mobilnät.</span><span class="sxs-lookup"><span data-stu-id="bd687-131">After updating, it executes two queries: hello first selects only hello device twins of devices located in hello **Redmond43** plant, and hello second refines hello query tooselect only hello devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="bd687-132">Observera att hello föregående kod när den skapar hello **frågan** objekt, anger ett maximalt antal returnerade dokument.</span><span class="sxs-lookup"><span data-stu-id="bd687-132">Note that hello previous code, when it creates hello **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="bd687-133">Hej **frågan** objektet innehåller en **HasMoreResults** boolesk egenskap som du kan använda tooinvoke hello **GetNextAsTwinAsync** metoder flera gånger tooretrieve alla resultat.</span><span class="sxs-lookup"><span data-stu-id="bd687-133">hello **query** object contains a **HasMoreResults** boolean property that you can use tooinvoke hello **GetNextAsTwinAsync** methods multiple times tooretrieve all results.</span></span> <span data-ttu-id="bd687-134">En metod som kallas **GetNextAsJson** är tillgänglig för resultat som är inte enheten twins, till exempel resultatet av aggregering frågor.</span><span class="sxs-lookup"><span data-stu-id="bd687-134">A method called **GetNextAsJson** is available for results that are not device twins, for example, results of aggregation queries.</span></span>
1. <span data-ttu-id="bd687-135">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="bd687-135">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. <span data-ttu-id="bd687-136">Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **AddTagsAndQuery** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="bd687-136">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **AddTagsAndQuery** project is **Start**.</span></span> <span data-ttu-id="bd687-137">Skapa hello lösning.</span><span class="sxs-lookup"><span data-stu-id="bd687-137">Build hello solution.</span></span>
1. <span data-ttu-id="bd687-138">Kör det här programmet genom att högerklicka på hello **AddTagsAndQuery** projekt och välja **felsöka**, följt av **Starta ny instans**.</span><span class="sxs-lookup"><span data-stu-id="bd687-138">Run this application by right-clicking on hello **AddTagsAndQuery** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="bd687-139">Du bör se en enhet i hello resultat för hello frågan ställa för alla enheter i **Redmond43** och ingen för hello-fråga som begränsar hello resultatet toodevices som använder ett mobilnät.</span><span class="sxs-lookup"><span data-stu-id="bd687-139">You should see one device in hello results for hello query asking for all devices located in **Redmond43** and none for hello query that restricts hello results toodevices that use a cellular network.</span></span>
   
    ![Resultatet av frågan i fönstret][img-addtagapp]

<span data-ttu-id="bd687-141">I nästa avsnitt om hello, skapar du en enhetsapp som rapporterar hello anslutningsinformation och ändringar hello resultatet av frågan hello hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bd687-141">In hello next section, you create a device app that reports hello connectivity information and changes hello result of hello query in hello previous section.</span></span>

## <a name="create-hello-device-app"></a><span data-ttu-id="bd687-142">Skapa hello enhetsapp</span><span class="sxs-lookup"><span data-stu-id="bd687-142">Create hello device app</span></span>
<span data-ttu-id="bd687-143">I det här avsnittet skapar du en Node.js-konsolprogram som ansluter tooyour hubb som **myDeviceId**, och sedan uppdaterar rapporterade egenskaper toocontain hello informationen att servern är ansluten med hjälp av ett mobilnät.</span><span class="sxs-lookup"><span data-stu-id="bd687-143">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, and then updates its reported properties toocontain hello information that it is connected using a cellular network.</span></span>

1. <span data-ttu-id="bd687-144">Skapa en ny tom mapp som kallas **reportconnectivity**.</span><span class="sxs-lookup"><span data-stu-id="bd687-144">Create a new empty folder called **reportconnectivity**.</span></span> <span data-ttu-id="bd687-145">I hello **reportconnectivity** mapp, skapa en ny package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="bd687-145">In hello **reportconnectivity** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="bd687-146">Acceptera alla standardvärden för hello.</span><span class="sxs-lookup"><span data-stu-id="bd687-146">Accept all hello defaults.</span></span>
   
    ```
    npm init
    ```
1. <span data-ttu-id="bd687-147">Vid en kommandotolk i hello **reportconnectivity** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet**, och **azure-iot-enhet – mqtt** paketet :</span><span class="sxs-lookup"><span data-stu-id="bd687-147">At your command prompt in hello **reportconnectivity** folder, run hello following command tooinstall hello **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. <span data-ttu-id="bd687-148">Med hjälp av en textredigerare, skapa en ny **ReportConnectivity.js** filen i hello **reportconnectivity** mapp.</span><span class="sxs-lookup"><span data-stu-id="bd687-148">Using a text editor, create a new **ReportConnectivity.js** file in hello **reportconnectivity** folder.</span></span>
1. <span data-ttu-id="bd687-149">Lägg till följande kod toohello hello **ReportConnectivity.js** filen och ersätta hello platshållare för enheten anslutningssträngen med hello något du kopierade när du skapade hello **myDeviceId** enhet identitet:</span><span class="sxs-lookup"><span data-stu-id="bd687-149">Add hello following code toohello **ReportConnectivity.js** file, and substitute hello placeholder for device connection string with hello one you copied when you created hello **myDeviceId** device identity:</span></span>
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
   
            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };
   
                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
   
    <span data-ttu-id="bd687-150">Hej **klienten** objektet innehåller alla hello-metoder som du behöver toointeract med enheten twins från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="bd687-150">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="bd687-151">Hej föregående kod när den initierar hello **klienten** objekt, hämtar hello enheten dubbla för **myDeviceId** och uppdaterar egenskapen rapporterade med hello anslutningsinformation.</span><span class="sxs-lookup"><span data-stu-id="bd687-151">hello previous code, after it initializes hello **Client** object, retrieves hello device twin for **myDeviceId** and updates its reported property with hello connectivity information.</span></span>
1. <span data-ttu-id="bd687-152">Kör hello enhetsapp</span><span class="sxs-lookup"><span data-stu-id="bd687-152">Run hello device app</span></span>
   
        node ReportConnectivity.js
   
    <span data-ttu-id="bd687-153">Du bör se hello-meddelande `twin state reported`.</span><span class="sxs-lookup"><span data-stu-id="bd687-153">You should see hello message `twin state reported`.</span></span>
1. <span data-ttu-id="bd687-154">Nu när hello enheten rapporterade dess anslutningsinformation, bör den visas i båda frågor.</span><span class="sxs-lookup"><span data-stu-id="bd687-154">Now that hello device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="bd687-155">Kör hello .NET **AddTagsAndQuery** app toorun hello frågar igen.</span><span class="sxs-lookup"><span data-stu-id="bd687-155">Run hello .NET **AddTagsAndQuery** app toorun hello queries again.</span></span> <span data-ttu-id="bd687-156">Den här gången **myDeviceId** ska visas i båda frågeresultaten.</span><span class="sxs-lookup"><span data-stu-id="bd687-156">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![][img-addtagapp2]

## <a name="next-steps"></a><span data-ttu-id="bd687-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd687-157">Next steps</span></span>
<span data-ttu-id="bd687-158">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="bd687-158">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="bd687-159">Du lagt till enhetsmetadata som taggar från en backend-app och skrev en simulerad enhet app tooreport anslutning enhetsinformation i hello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="bd687-159">You added device metadata as tags from a back-end app, and wrote a simulated device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="bd687-160">Du också lära sig hur tooquery informationen med hjälp av frågespråket för hello SQL-liknande IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bd687-160">You also learned how tooquery this information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="bd687-161">Använd hello följande resurser toolearn hur till:</span><span class="sxs-lookup"><span data-stu-id="bd687-161">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="bd687-162">Skicka telemetri från enheter med hello [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="bd687-162">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="bd687-163">Konfigurera enheter med hjälp av enheten två egenskaper med hello [Använd önskad egenskaper tooconfigure enheter] [ lnk-twin-how-to-configure] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="bd687-163">configure devices using device twin's desired properties with hello [Use desired properties tooconfigure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="bd687-164">Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app) med hello [metoder från direkt] [ lnk-methods-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="bd687-164">control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

